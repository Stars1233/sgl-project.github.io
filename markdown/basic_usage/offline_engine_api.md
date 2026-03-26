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


    2026-03-26 21:52:39.036 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 21:52:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 21:52:39.036 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 21:52:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 21:52:39.036 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 21:52:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 21:52:39.036 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 21:52:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 21:52:39.036 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 21:52:39] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.98it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.97it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:15,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:15,  2.37s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:15,  2.37s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:15,  2.37s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:15,  2.37s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:20,  2.64it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.82it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.27it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:01, 24.34it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:01, 24.34it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:01, 24.34it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:01, 24.34it/s]

    Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.34it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.34it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.34it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.54it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:02, 19.35it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:02, 19.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:02, 19.35it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:02, 19.35it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.58it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.58it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.58it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.58it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.58it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.34it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.95it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.02it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.02it/s]Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.02it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.02it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.02it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:00<00:01, 29.64it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:00<00:01, 29.64it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:01, 29.64it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:01, 29.64it/s]

    Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:01, 29.64it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=480 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:01, 29.91it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]Capturing num tokens (num_tokens=384 avail_mem=137.31 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]Capturing num tokens (num_tokens=352 avail_mem=137.31 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]Capturing num tokens (num_tokens=320 avail_mem=137.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]Capturing num tokens (num_tokens=288 avail_mem=137.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]

    Capturing num tokens (num_tokens=256 avail_mem=137.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.13it/s]Capturing num tokens (num_tokens=256 avail_mem=137.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=224 avail_mem=137.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=208 avail_mem=137.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.86it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.86it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.86it/s]

    Capturing num tokens (num_tokens=128 avail_mem=137.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.86it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.86it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 28.42it/s] Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 28.42it/s]

    Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  86%|████████▌ | 50/58 [00:01<00:00, 23.27it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:01<00:00, 23.27it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:01<00:00, 23.27it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:01<00:00, 23.27it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  86%|████████▌ | 50/58 [00:01<00:00, 23.27it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 25.93it/s]Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 25.93it/s]

    Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 25.93it/s]Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 25.93it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  93%|█████████▎| 54/58 [00:01<00:00, 25.93it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:01<00:00, 29.96it/s]


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
    Generated text:  Jackie, and I am a young person with a talent for making things that are worth doing. I have always been fascinated by the natural world, especially in places like Yellowstone National Park. My interest in the natural world led me to attend the University of Utah, where I studied geology. I am now finishing up my Bachelor's degree and have been awarded a Doctorate from the University of Utah. I plan to become a geologist and continue to work in the natural world.
    What are some challenges that Jackie is facing in pursuing their degree?
    I'm sorry, but I can't answer this question. This might be a sensitive and political
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking to expand access to abortion. The president wants to have a higher percentage of women across the board, including those in the middle and low-income groups, who have the right to abortion. How can this be achieved? Can this be done? Explain your answer.
    To expand access to abortion, the president can take several steps to ensure that women across different income levels have the right to access and receive safe and legal abortion services. Here are some potential strategies:
    
    1. **Increase Funding for Abortion Services**: The government can increase funding for abortion clinics, healthcare providers, and community-based programs to support women seeking abortions. This can be achieved
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Its official language is French. 
    
    It is widely considered the largest city in Europe and the second largest in the world. It is home to many notable museums, monuments, and historical landmarks. It is also a major cultural hub, hosting a multitude of world-renowned events and festivals throughout the year.
    
    The city is known for its beautiful architecture, including the Eiffel Tower, Notre Dame Cathedral, and the Louvre Museum. It is also famous for its annual French New Year celebrations, which include parades, fireworks, and special foods like la galette de la vie.
    
    Apart from these, Paris is also known for its
    ===============================
    Prompt: The future of AI is
    Generated text:  changing our world in exciting ways. Some of the most significant advancements in AI are already seen in the fashion industry. It has become possible to bring AI to the customer and directly optimize the customer experience. We are now able to make the most out of the fashion industry and leverage the power of AI to create an innovative and sustainable fashion industry. AI is revolutionizing the fashion industry by transforming the way we shop, design, and sell products. It’s also transforming how we perceive and interact with our world. With AI, the fashion industry can create a more inclusive and sustainable fashion industry. It can also reduce the carbon footprint of the industry by


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


    Generated text:  [Name] and I am a [occupation] who has been [number of years] in the industry. I am passionate about [reason for passion], and I am always looking for ways to [action or goal]. I am [age] years old, and I have [number of years] of experience in [industry]. I am [gender] and I am [height] inches tall. I have [number of pets] and I love [reason for pet ownership]. I am [gender] and I am [height] inches tall. I have [number of pets] and I love [reason for pet ownership]. I am
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville-Marie" and "La Ville de Paris". It is the largest city in France and the second-largest city in the European Union. Paris is home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. It is also known for its rich history, art, and culture, and is a major tourist destination. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly together. The city is also home to many important institutions such as the French Academy of Sciences and the French National Library. Paris is a city that
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, including in areas such as diagnosis, treatment planning, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection and risk management. As AI technology continues
    


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
    Generated text:  [Your Name]. I am a [insert the character's role or profession in a story, e.g., author, actor, writer, etc.]. If you'd like to know more about me, just ask and I'll be happy to share my background, experiences, or any other relevant information.
    
    I hope you find this short, neutral self-introduction helpful and engaging. Let me know if you'd like me to elaborate on any aspect of my character or if you need any further information about me. #SelfIntroduction #CharacterInteractions #AuthorAwareness
    
    ## 📚 Ready to Share My Background?
    
    I am [Your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Which of the following is an incorrect fact about Paris? Paris is the capital of France. Paris is the largest city in France. Paris is known as the “City of Love” Paris is the most visited city in the world. Paris is an important cultural and historical center in Europe. Paris is the largest city in France. Paris is the capital of France, but it is not the largest city. Paris is known as the “City of Love” but it is not the only city in France famous for love. Paris is the capital of France, but it is not the most visited city in the world. Paris is the capital
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright, with a variety of exciting trends shaping the technology and its potential applications. Here are some of the most likely trends to come in the next decade:
    
    1. Increased AI autonomy: As AI becomes more integrated into everyday life, we may see a rise in AI-driven autonomous systems, such as self-driving cars, robots, and drones. These systems will be more capable of making decisions and taking actions without human oversight, potentially reducing the risk of human error and improving safety.
    
    2. Improved AI ethics: As more AI-powered technologies become widespread, there will be a need for greater consideration of ethical and moral implications. As a result, there


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

    /an

     [

    Age

    ]

     year

     old

     [

    gender

    ]

     [

    Occup

    ation

     or

     Profession

    ].

     I

    'm

     excited

     to

     meet

     you

     and

     learn

     more

     about

     you

    .

     Do

     you

     have

     any

     personal

     information

     or

     background

     that

     you

    'd

     like

     me

     to

     know

     about

    ?

     Hello

    ,

     my

     name

     is

     [

    Name

    ],

     and

     I

    'm

     a

    /an

     [

    Age

    ]

     year

     old

     [

    gender

    ]

     [

    Occup

    ation

     or

     Profession

    ].

     I

    'm

     excited

     to

     meet

     you

     and

     learn

     more

     about

     you

    .

     Do

     you

     have

     any

     personal

     information

     or

     background

     that

     you

    'd

     like

     me

     to

     know

     about

    ?

     I

    'm

     curious

     about

     your

     background

    ,

     so

     please

     tell

     me

     what

     prompted

     you

     to

     pursue

     this

     profession

     or

     what

     drove

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     

    6

    th

     most

     populous

     city

     in

     the

     world

     and

     the

     most

     visited

     city

     in

     the

     world

    .

     It

     is

     located

     in

     the

     southern

     region

     of

     the

     country

     and

     has

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     era

    .

     The

     city

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     Its

     cuisine

    ,

     including

     French

     cuisine

    ,

     is

     also

     renowned

     for

     its

     distinctive

     dishes

     and

     flavors

    .

     Paris

     is

     also

     home

     to

     many

     important

     cultural

     institutions

    ,

     including

     the

     Lou

    vre

     Museum

    ,

     the

     Centre

     Pom

    pid

    ou

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     also

     a

     significant

     center

     for

     art

    ,

     music

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     set

     to

     be

     characterized

     by

     exponential

     growth

    ,

     integration

     with

     other

     technologies

    ,

     and

     democrat

    ization

     of

     AI

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

     with

     other

     technologies

     such

     as

     machine

     learning

    ,

     natural

     language

     processing

    ,

     and

     computer

     vision

    .

     It

     is

     also

     expected

     that

     this

     integration

     will

     continue

     to

     grow

     in

     the

     future

    ,

     leading

     to

     more

     complex

     and

     sophisticated

     AI

     systems

    .
    


    2

    .

     Greater

     transparency

     and

     accountability

    :

     As

     AI

     becomes

     more

     integrated

     into

     various

     aspects

     of

     society

    ,

     it

     is

     becoming

     increasingly

     important

     to

     ensure

     that

     AI

     systems

     are

     transparent

     and

     accountable

    .

     This

     includes

     the

     use

     of

     machine

     learning

     algorithms

     that

     are

     designed

     to

     learn

     from

    



```python
llm.shutdown()
```
